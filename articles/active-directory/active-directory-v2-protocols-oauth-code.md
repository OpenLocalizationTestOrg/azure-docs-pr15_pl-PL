

<properties
    pageTitle="Azure AD 2.0 OAuth autoryzacji kod przepływu | Microsoft Azure"
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
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>Protokoły 2.0 — przepływ kod autoryzacji OAuth 2.0

Udzielanie kodu autoryzacji OAuth 2.0 można używać w aplikacji, które są zainstalowane na urządzeniu, uzyskanie dostępu do chronionych zasoby, takie jak interfejsy API sieci web.  Przy użyciu aplikacji 2.0 modelu stosowania OAuth 2.0, możesz dodać logowanie się i interfejsu API dostępu do swoich urządzeń przenośnych i stacjonarnych aplikacji.  Ten przewodnik jest niezależny od języka i opisano, jak wysyłać i odbierać wiadomości HTTP bez przy użyciu jednej z naszych bibliotek Otwórz źródło.

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

Przepływ kod autoryzacji OAuth 2.0 opis znajduje się w [sekcji 4.1 specyfikacji OAuth 2.0](http://tools.ietf.org/html/rfc6749).  Służy do wykonywania uwierzytelniania i autoryzacji w większości typów aplikacji, takich jak [aplikacje sieci web](active-directory-v2-flows.md#web-apps) i [oryginalnie zainstalowane aplikacje](active-directory-v2-flows.md#mobile-and-native-apps).  Umożliwia aplikacji bezpieczne uzyskanie access_tokens, dzięki czemu można uzyskać dostęp do zasobów, które są chronione za pomocą punkt końcowy 2.0.  

## <a name="protocol-diagram"></a>Diagram Protocol (protokół)
Na wysokim poziomie przepływ całego uwierzytelniania dla aplikacji macierzystego mobile wygląda nieco następująco:

![Przepływ uwierzytelniania kod OAuth](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Żądanie kodu autoryzacji
Przepływ kodu autoryzacji zaczyna się od klienta kierujące użytkownikowi `/authorize` punktu końcowego.  W tym żądaniu klienta wskazuje uprawnienia, które trzeba ją uzyskać od użytkownika:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] Kliknij łącze poniżej, aby wykonać to żądanie! Po zalogowaniu się, przeglądarki, nastąpi przekierowanie do `https://localhost/myapp/` z `code` na pasku adresu.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>

| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| dzierżawy | Wymagane | `{tenant}` Wartość w polu Ścieżka żądania może być używana w celu kontrolowania, kto może zalogować się do aplikacji.  Są dozwolone wartości `common`, `organizations`, `consumers`i dzierżawa identyfikatory.  Aby uzyskać więcej szczegółów zobacz [podstawy Protocol (protokół)](active-directory-v2-protocols.md#endpoints). |
| client_id | Wymagane | Identyfikator aplikacji przypisania aplikacji portalu rejestracji ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)). |
| response_type | Wymagane | Musi zawierać `code` przepływ kodu autoryzacji. |
| redirect_uri | zalecane | Redirect_uri aplikacji, której uwierzytelniania odpowiedzi można wysyłane i otrzymywane przez aplikację.  Dokładnie musi odpowiadać jeden z redirect_uris, które zarejestrowane w portalu, z wyjątkiem musi być zakodowany w adresie url.  W przypadku aplikacji natywne i urządzeń przenośnych, należy używać wartości domyślnej `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| zakres | Wymagane | Rozdzielany spacjami [zakresy](active-directory-v2-scopes.md) , które ma użytkownik wyraża zgodę na.  |
| response_mode | zalecane | Określa metodę, których należy używać wysyłanie wyniku wstecz token do aplikacji.  Może być `query` lub `form_post`.  |
| Województwo | zalecane | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token.  Może być ciągiem dowolną zawartość, która ma.  Losowo wygenerowanym wartości unikatowe jest zazwyczaj używany służące do [zapobiegania powstawaniu atakami fałszowaniu żądanie między witrynami](http://tools.ietf.org/html/rfc6749#section-10.12).  Stan służy także do kodowania informacji o stanie użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony lub widoku, w których znajdowały się na. |
| wiersz | opcjonalne | Wskazuje typ interakcji użytkownika, który jest wymagane.  Jedyne prawidłowe wartości w tej chwili to "logowania", "Brak" i "zgoda".  `prompt=login`będzie wymusić na użytkowniku wprowadź swoje poświadczenia na wniosku, Negacja logowania jednokrotnego na.  `prompt=none`odwrotny - go będziesz mieć pewność, że użytkownik nie jest wyświetlany wszystkie interaktywne monity jakiejkolwiek.  Jeśli żądanie nie może się zakończyć w trybie cichym za pośrednictwem logowania jednokrotnego na, punkt końcowy 2.0 — powoduje zwrócenie błędu.  `prompt=consent`wywoła okno dialogowe zgody OAuth, gdy użytkownik zaloguje, użytkownikowi uprawnienia do aplikacji. |
| login_hint | opcjonalne | Można wstępnie wypełnianie pola Adres nazwa_użytkownika/e-mail stronie logowania dla użytkownika, jeśli znasz jego nazwę użytkownika wyprzedzeniem.  Aplikacje często użyje ten parametr podczas ponownego uwierzytelniania już masz wyodrębnionych nazwa użytkownika z poprzedniego logowania przy użyciu `preferred_username` rościć sobie. |
| domain_hint | opcjonalne | Może być jednym z `consumers` lub `organizations`.  Jeśli zawiera, pominie procesu odnajdowania opartej na poczcie e-mail tego użytkownika przez moduł na stronie logowania 2.0, prowadzące do nieco usprawnionym środowisko użytkownika.  Aplikacje często użyje ten parametr podczas ponownego uwierzytelniania po wyodrębnieniu `tid` z poprzedniego logowania.  Jeśli `tid` rościć sobie wartość jest `9188040d-6c67-4c5b-b112-36a304b66dad`, należy użyć `domain_hint=consumers`.  W przeciwnym razie za pomocą `domain_hint=organizations`. |

Na tym etapie użytkownik zostanie poproszony wprowadź swoje poświadczenia i ukończyć uwierzytelniania.  Punkt końcowy 2.0 zapewni również, że użytkownik ma zgodę na uprawnienia określone w `scope` kwerenda parametryczna.  Jeśli użytkownik nie ma zgodę na dowolny z tych uprawnień, zostanie wyświetlone pytanie użytkownikowi wyraża zgodę na wymagane uprawnienia.  Szczegóły dotyczące [uprawnień, zgoda oraz wielu dzierżawy aplikacji znajdują się w tym miejscu](active-directory-v2-scopes.md).

Gdy użytkownik uwierzytelnia i udziela zgody, punkt końcowy 2.0 zwróci odpowiedź dotycząca aplikacji na wskazanej `redirect_uri`, przy użyciu metody określonej w `response_mode` parametru.

#### <a name="successful-response"></a>Pomyślną odpowiedź
Pomyślną odpowiedź przy użyciu `response_mode=query` wygląda jak:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Kod | Authorization_code, tej aplikacji. Aplikacja umożliwia kodu autoryzacji żądanie token dostępu dla zasobu docelowego.  Authorization_codes są bardzo krótko, zwykle wygasają po około 10 minut. |
| Województwo | Jeśli parametr Stan znajduje się w wezwaniu na tej samej wartości powinien być wyświetlany w odpowiedzi. Aplikacja należy sprawdzić, czy wartości stanu na żądanie i odpowiedź są identyczne. |

#### <a name="error-response"></a>Odpowiedzi komunikat o błędzie
Odpowiedzi na błędy mogą być również wysyłane do `redirect_uri` , aplikacja może podejmie wobec nich odpowiednio:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kodu błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Kody błędów autoryzacji punktu końcowego błędów

W poniższej tabeli opisano różne kody błędów, które mogą zostać zwrócone w `error` parametru odpowiedzi błędu.

| Kod błędu | Opis | Akcja klienta |
|------------|-------------|---------------|
| invalid_request | Błąd protokołu, taki jak brak wymaganego parametru. | Rozwiąż i prześlij żądanie. To jest rozwinięcie błąd jest zwykle otrzymanych podczas testowania początkowej.|
| unauthorized_client | Aby zażądać kod autoryzacji nie jest dozwolona z aplikacją kliencką. | Zwykle dzieje się tak, gdy aplikacja klienta nie jest zarejestrowany w Azure AD lub nie są dodawane do dzierżawy Azure AD użytkownika. Aplikacja może Monituj o instrukcje dotyczące instalowania aplikacji i dodanie go do Azure AD. |
| ACCESS_DENIED | Odmowa zgody właściciela zasobu | Z aplikacją kliencką może powiadomić użytkownika, którego nie można kontynuować, chyba że użytkownik wyraża zgodę. |
| unsupported_response_type | W wezwaniu na serwerze autoryzacji nie obsługuje typu odpowiedzi. | Rozwiąż i prześlij żądanie. To jest rozwinięcie błąd jest zwykle otrzymanych podczas testowania początkowej.|
|server_error | Serwer wystąpił nieoczekiwany błąd. | Ponawianie żądania. Tych błędów może spowodować tymczasowe warunków. Z aplikacją kliencką mogą wyjaśnić użytkownikowi, że odpowiedzi jest opóźnione ukończenia tymczasowy błąd. |
| temporarily_unavailable | Serwer jest tymczasowo zajęta do obsługi żądania. | Ponawianie żądania. Z aplikacją kliencką może wyjaśniono użytkownikowi, że odpowiedzi jest opóźnione ukończenia tymczasowe warunku. |
| invalid_resource |Zasobu docelowego jest nieprawidłowy, ponieważ nie istnieje, nie można go znaleźć Azure AD lub nie jest poprawnie skonfigurowany.| Oznacza to, że zasobu, jeśli istnieje, nie został skonfigurowany w dzierżawie. Aplikacja może Monituj o instrukcje dotyczące instalowania aplikacji i dodanie go do Azure AD. |

## <a name="request-an-access-token"></a>Żądanie token dostępu
Teraz, gdy zostało nabyte authorization_code i udzielono uprawnień przez użytkownika, można zrealizować `code` dla `access_token` do odpowiedniej zasobu, wysyłając `POST` żądanie `/token` punkt końcowy:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Wypróbuj wykonywania tego żądania w Postman! (Pamiętaj zamienić `code`)     [ ![Uruchamiane w Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------------- |
| dzierżawy | Wymagane | `{tenant}` Wartość w polu Ścieżka żądania może być używana w celu kontrolowania, kto może zalogować się do aplikacji.  Są dozwolone wartości `common`, `organizations`, `consumers`i dzierżawa identyfikatory.  Aby uzyskać więcej szczegółów zobacz [podstawy Protocol (protokół)](active-directory-v2-protocols.md#endpoints). |
| client_id | Wymagane | Identyfikator aplikacji przypisania aplikacji portalu rejestracji ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)). |
| grant_type | Wymagane | Musi być `authorization_code` dla przepływu kodu autoryzacji. |
| zakres | Wymagane | Rozdzielany spacjami zakresów.  Zakresy w tym nogi musi być równoważne lub podzbiór zakresów wymagane w pierwszym nogi.  Jeśli zakresy określone w tym żądaniu obejmują wiele serwerów zasobów, punkt końcowy 2.0 zwróci tokenu dla zasobu określonych w zakresie pierwszego.  Bardziej szczegółowy opis zakresów można znaleźć [uprawnienia, zgoda oraz zakresów](active-directory-v2-scopes.md).  |
| Kod | Wymagane | Authorization_code, które zostało nabyte w pierwszym nogi przepływu.   |
| redirect_uri | Wymagane | Tę samą liczbę redirect_uri, który został użyty do uzyskania authorization_code. |
| client_secret | wymagane dla aplikacji sieci web | Hasło aplikacji utworzonej w portalu rejestracji aplikacji dla aplikacji.  Nie należy można użyć w natywnej aplikacji, ponieważ client_secrets nie mogą być w wiarygodny sposób przechowywane na urządzeniach.  Jest wymagane dla aplikacji sieci web i interfejsów API, które mają możliwość bezpieczne przechowywanie client_secret po stronie serwera w sieci web. |

#### <a name="successful-response"></a>Pomyślną odpowiedź
Pomyślną odpowiedź token będzie wyglądać:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametr | Opis |
| ----------------------- | ------------------------------- |
| access_token | Token dostępu wymagane. Aplikacja umożliwia ten token uwierzytelniania zabezpieczonego zasobu, takiego jak interfejsu API sieci web. |
| token_type | Wskazuje typ tokenu wartość. Tylko typu, która obsługuje Azure AD jest okaziciela  |
| expires_in | Jak długo trwa token dostępu jest prawidłowy (w sekundach). |
| zakres | Zakresy, które access_token dotyczy. |
| refresh_token |  Token OAuth 2.0 odświeżania. Aplikacja służy ten token uzyskiwanie tokeny dostępu dodatkowe po wygaśnięciu bieżącego tokenu dostępu.  Refresh_tokens są długotrwałe i pozwalają zachować dostęp do zasobów przez dłuższy czas.  Aby uzyskać więcej szczegółów zapoznaj się z [odwołania do tokenu 2.0](active-directory-v2-tokens.md).  |
| id_token | Niepodpisany JSON Web Token (JWT). Base64Url mogą aplikacji dekodowanie segmenty ten token na żądanie informacji o użytkownik zalogowany. Aplikacja może pamięci podręcznej wartości i wyświetlać je, ale go nie będą miały od nich autoryzacji lub ograniczenia zabezpieczeń.  Aby uzyskać więcej informacji na temat id_tokens zobacz [odwołania do tokenu końcowego 2.0](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Odpowiedzi komunikat o błędzie
Błąd odpowiedzi będzie wyglądać:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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
| Błąd | Ciąg kodu błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania.  |
| error_codes | Lista kodów błędów określone usługi STS, które mogą ułatwić diagnostyki.  |
| Sygnatura czasowa | Czas, w którym wystąpił błąd. |
| trace_id | Unikatowy identyfikator żądania, które mogą być pomocne w diagnostyczne.  |
| correlation_id | Unikatowy identyfikator żądania, które mogą być pomocne w diagnostyki elementów. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Kody błędów pod kątem błędów token punktu końcowego

| Kod błędu | Opis | Akcja klienta |
|------------|-------------|---------------|
| invalid_request | Błąd protokołu, taki jak brak wymaganego parametru. | Naprawianie i prześlij żądanie |
| invalid_grant | Kod autoryzacji jest nieprawidłowa lub wygasła. | Spróbuj nowe żądanie do `/authorize` punktu końcowego |
| unauthorized_client | Aby użyć tego typu Udziel autoryzacji nie jest autoryzowany uwierzytelniony klient. | Zwykle dzieje się tak, gdy aplikacja klienta nie jest zarejestrowany w Azure AD lub nie są dodawane do dzierżawy Azure AD użytkownika. Aplikacja może Monituj o instrukcje dotyczące instalowania aplikacji i dodanie go do Azure AD. |
| invalid_client | Uwierzytelnianie klienta na podstawie nie powiodło się. | Poświadczenia klienta są nieprawidłowe. Aby rozwiązać problem, administrator aplikacji aktualizuje poświadczenia. |
| unsupported_grant_type | Serwer autoryzacji nie obsługuje typ Udziel autoryzacji. | Zmienianie typu udzielanie w wezwaniu. Ten typ błędu powinna zostać wykonana tylko w czasie projektowania i wykrywania podczas testowania początkowej. |
| invalid_resource | Zasobu docelowego jest nieprawidłowy, ponieważ nie istnieje, nie można go znaleźć Azure AD lub nie jest poprawnie skonfigurowany. | Oznacza to, że zasobu, jeśli istnieje, nie został skonfigurowany w dzierżawie. Aplikacja może Monituj o instrukcje dotyczące instalowania aplikacji i dodanie go do Azure AD. |
| interaction_required | Żądanie wymaga interakcji użytkownika. Na przykład krok dodatkowego uwierzytelniania jest wymagany. | Ponawianie żądania z tego zasobu. |
| temporarily_unavailable | Serwer jest tymczasowo zajęta do obsługi żądania. | Ponawianie żądania. Z aplikacją kliencką może wyjaśniono użytkownikowi, że odpowiedzi jest opóźnione ukończenia tymczasowe warunku.|

## <a name="use-the-access-token"></a>Używanie token dostępu
Teraz, gdy został pomyślnie pozyskano `access_token`, można używać tokenu w żądania do interfejsów API usługi sieci Web, w tym `Authorization` nagłówka:

> [AZURE.TIP] Wykonanie tego żądania w Postman! (Zamień `Authorization` nagłówek pierwszej)     [ ![Uruchamiane w Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Odświeżanie token dostępu
Access_tokens są krótkie żyła i należy je odświeżyć, po ich wygaśnięciu, aby kontynuować, uzyskiwanie dostępu do zasobów.  Możesz to zrobić przesyłając innego `POST` żądanie `/token` punktu końcowego, tym razem zapewniające `refresh_token` zamiast `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Wypróbuj wykonywania tego żądania w Postman! (Pamiętaj zamienić `refresh_token`)     [ ![Uruchamiane w Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parametr | | Opis |
| ----------------------- | ------------------------------- | -------- |
| dzierżawy | Wymagane | `{tenant}` Wartość w polu Ścieżka żądania może być używana w celu kontrolowania, kto może zalogować się do aplikacji.  Są dozwolone wartości `common`, `organizations`, `consumers`i dzierżawa identyfikatory.  Aby uzyskać więcej szczegółów zobacz [podstawy Protocol (protokół)](active-directory-v2-protocols.md#endpoints). |
| client_id | Wymagane | Identyfikator aplikacji przypisania aplikacji portalu rejestracji ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)). |
| grant_type | Wymagane | Musi być `refresh_token` dla tego nogi przepływ kodu autoryzacji. |
| zakres | Wymagane | Rozdzielany spacjami zakresów.  Zakresy w tym nogi musi być równoważne lub podzbiór zakresów wymagane w oryginalnej nogi żądania authorization_code.  Jeśli zakresy określone w tym żądaniu obejmują wiele serwerów zasobów, punkt końcowy 2.0 zwróci tokenu dla zasobu określonych w zakresie pierwszego.  Bardziej szczegółowy opis zakresów można znaleźć [uprawnienia, zgoda oraz zakresów](active-directory-v2-scopes.md).  |
| refresh_token | Wymagane | Refresh_token, które zostało nabyte w drugim nogi przepływu.   |
| redirect_uri | Wymagane | Tę samą liczbę redirect_uri, który został użyty do uzyskania authorization_code. |
| client_secret | wymagane dla aplikacji sieci web | Hasło aplikacji utworzonej w portalu rejestracji aplikacji dla aplikacji.  Nie należy można użyć w natywnej aplikacji, ponieważ client_secrets nie mogą być w wiarygodny sposób przechowywane na urządzeniach.  Jest wymagane dla aplikacji sieci web i interfejsów API, które mają możliwość bezpieczne przechowywanie client_secret po stronie serwera w sieci web. |

#### <a name="successful-response"></a>Pomyślną odpowiedź
Pomyślną odpowiedź token będzie wyglądać:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametr | Opis |
| ----------------------- | ------------------------------- |
| access_token | Token dostępu wymagane. Aplikacja umożliwia ten token uwierzytelniania zabezpieczonego zasobu, takiego jak interfejsu API sieci web. |
| token_type | Wskazuje typ tokenu wartość. Tylko typu, która obsługuje Azure AD jest okaziciela  |
| expires_in | Jak długo trwa token dostępu jest prawidłowy (w sekundach). |
| zakres | Zakresy, które access_token dotyczy. |
| refresh_token |  Nowy token odświeżania OAuth 2.0. Stare token odświeżania należy zastąpić token ten nowo pobranych Odśwież, aby upewnić się, że usługi tokeny odświeżania ważność tak długo.  |
| id_token | Niepodpisany JSON Web Token (JWT). Base64Url mogą aplikacji dekodowanie segmenty ten token na żądanie informacji o użytkownik zalogowany. Aplikacja może pamięci podręcznej wartości i wyświetlać je, ale go nie będą miały od nich autoryzacji lub ograniczenia zabezpieczeń.  Aby uzyskać więcej informacji na temat id_tokens zobacz [odwołania do tokenu końcowego 2.0](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Odpowiedzi komunikat o błędzie
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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
| Błąd | Ciąg kodu błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania.  |
| error_codes | Lista kodów błędów określone usługi STS, które mogą ułatwić diagnostyki.  |
| Sygnatura czasowa | Czas, w którym wystąpił błąd. |
| trace_id | Unikatowy identyfikator żądania, które mogą być pomocne w diagnostyczne.  |
| correlation_id | Unikatowy identyfikator żądania, które mogą być pomocne w diagnostyki elementów. |

Opis akcji zalecane klienta i kody błędów zobacz [kody błędów pod kątem błędów token punktu końcowego](#error-codes-for-token-endpoint-errors).
