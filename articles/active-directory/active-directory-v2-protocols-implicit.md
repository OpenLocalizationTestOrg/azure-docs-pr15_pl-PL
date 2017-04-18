<properties
    pageTitle="Azure AD 2.0 niejawne przepływu | Microsoft Azure"
    description="Konstrukcyjnych aplikacji sieci web przy użyciu implementacji 2.0 Azure AD niejawne przepływu dla pojedynczej strony aplikacje."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---spas-using-the-implicit-flow"></a>2.0 protokoły - źródła przy użyciu niejawne przepływu
Z punktem końcowym 2.0 — użytkownicy mogą logowania się do aplikacji pojedynczej strony z kontami osobistych i służbowych/szkolnych od firmy Microsoft.  Pojedynczej strony i inne aplikacje JavaScript który Uruchom przede wszystkim na powierzchnię przeglądarki kilka interesujące wzywa, gdy nadejdzie uwierzytelnianie:

- Właściwości zabezpieczeń te aplikacje są znacznie różni się od tradycyjnych serwera aplikacji sieci web.
- Wiele serwery autoryzacji i dostawcy tożsamości nie obsługują żądania CORS.
- Cała strona przeglądarki przekierowuje od aplikacji stają się szczególnie inwazyjnych wrażenia użytkownika.

Dla tych aplikacji (Pomyśl: AngularJS, Ember.js, React.js, itp) Azure AD obsługuje przepływu udzielanie niejawne OAuth 2.0.  Niejawne przepływu opisano w [Specyfikacji 2.0 OAuth](http://tools.ietf.org/html/rfc6749#section-4.2).  Podstawowy korzyści jest możliwość aplikację, aby uzyskać tokenów z Azure AD bez wykonywania serwera wewnętrznej bazy danych poświadczeń programu exchange.  Dzięki temu aplikację, aby zalogować się użytkownik, obsługa sesji i uzyskać tokenów innych sieci Web interfejsy API w kliencie kodu JavaScript.  Istnieje kilka ważne zagadnienia dotyczące zabezpieczeń wziąć pod uwagę podczas używania niejawnych przepływu - specjalnie wokół [klienta](http://tools.ietf.org/html/rfc6749#section-10.3) i [personifikacji użytkownika](http://tools.ietf.org/html/rfc6749#section-10.3).

Jeśli chcesz dodać uwierzytelniania do aplikacji JavaScript przy użyciu przepływu niejawne i Azure AD, zalecamy zapoznanie używasz naszych biblioteka kodu JavaScript Otwórz źródło [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  Istnieje kilka samouczki AngularJS [tutaj](active-directory-appmodel-v2-overview.md#getting-started) ułatwiające rozpoczęcie pracy.  

Jednak jeśli wolisz nie używać biblioteki w aplikacji pojedynczej strony i wysyłanie wiadomości protokołu, samodzielnie, wykonaj poniższe czynności ogólne.

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).
    
## <a name="protocol-diagram"></a>Diagram Protocol (protokół)
Niejawne całego Zaloguj przepływu wygląda mniej więcej tak — poszczególne kroki opisane szczegółowo poniżej.

![Łączenie OpenId torów](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Wyślij żądanie logowania

Początkowo logowania użytkownika do aplikacji, możesz wysłać żądanie autoryzacji [Łączenie OpenID](active-directory-v2-protocols-oidc.md) i uzyskać `id_token` od punktu końcowego 2.0:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [AZURE.TIP] Kliknij łącze poniżej, aby wykonać to żądanie! Po zalogowaniu się, przeglądarki, nastąpi przekierowanie do `https://localhost/myapp/` z `id_token` na pasku adresu.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>

| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| dzierżawy | Wymagane | `{tenant}` Wartość w polu Ścieżka żądania może być używana w celu kontrolowania, kto może zalogować się do aplikacji.  Są dozwolone wartości `common`, `organizations`, `consumers`i dzierżawa identyfikatory.  Aby uzyskać więcej szczegółów zobacz [podstawy Protocol (protokół)](active-directory-v2-protocols.md#endpoints). |
| client_id | Wymagane | Identyfikator aplikacji przypisania aplikacji portalu rejestracji ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)). |
| response_type | Wymagane | Musi zawierać `id_token` dla połączenia OpenID logowania.  Może też zawierać response_type `token`. Za pomocą `token` tutaj, aby umożliwić aplikacji do odbierania token dostępu od punktu końcowego Autoryzuj bez konieczności drugie żądanie do punktu końcowego Autoryzuj natychmiast.  Jeśli korzystasz z `token` response_type, `scope` parametr musi zawierać zakres wskazująca zasobów do wykonania token dla. |
| redirect_uri | zalecane | Redirect_uri aplikacji, której uwierzytelniania odpowiedzi można wysyłane i otrzymywane przez aplikację.  Dokładnie musi odpowiadać jeden z redirect_uris, które zarejestrowane w portalu, z wyjątkiem musi być zakodowany w adresie url. |
| zakres | Wymagane | Rozdzielany spacjami zakresów.  Dla OpenID połączyć, musi zawierać zakres `openid`, które przekłada się na uprawnienia "Zalogowanie użytkownika" w zgody interfejsu użytkownika.  Opcjonalnie możesz uwzględnienie `email` lub `profile` [zakresów](active-directory-v2-scopes.md) do uzyskania dostępu do dodatkowych danych.  Można także dodać inne zakresy w tym żądaniu żąda zgodę na różnych zasobów.  |
| response_mode | zalecane | Określa metodę, których należy używać wysyłanie wyniku wstecz token do aplikacji.  Należy `fragment` niejawne przepływu.  |
| Województwo | zalecane | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token.  Może być ciągiem dowolną zawartość, która ma.  Losowo wygenerowanym wartości unikatowe jest zazwyczaj używany służące do [zapobiegania powstawaniu atakami fałszowaniu żądanie między witrynami](http://tools.ietf.org/html/rfc6749#section-10.12).  Stan służy także do kodowania informacji o stanie użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony lub widoku, w których znajdowały się na. |
| Identyfikator jednorazowy | Wymagane | Wartość zawarte w wezwaniu na wygenerowane za pomocą aplikacji, które zostaną uwzględnione w wyniku id_token jako roszczenia.  Aplikacja może zweryfikować tę wartość na zmniejszeniu atakami token powtarzania.  Wartość jest zazwyczaj te, unikatowy ciąg, który może służyć do identyfikowania początku żądania.  |
| wiersz | opcjonalne | Wskazuje typ interakcji użytkownika, który jest wymagane.  Jedyne prawidłowe wartości w tej chwili to "logowania", "Brak" i "zgoda".  `prompt=login`będzie wymusić na użytkowniku wprowadź swoje poświadczenia na wniosku, Negacja logowania jednokrotnego na.  `prompt=none`odwrotny - go będziesz mieć pewność, że użytkownik nie jest wyświetlany wszystkie interaktywne monity jakiejkolwiek.  Jeśli żądanie nie może się zakończyć w trybie cichym za pośrednictwem logowania jednokrotnego na, punkt końcowy 2.0 — powoduje zwrócenie błędu.  `prompt=consent`wywoła okno dialogowe zgody OAuth, gdy użytkownik zaloguje, użytkownikowi uprawnienia do aplikacji. |
| login_hint | opcjonalne | Można wstępnie wypełnianie pola Adres nazwa_użytkownika/e-mail stronie logowania dla użytkownika, jeśli znasz jego nazwę użytkownika wyprzedzeniem.  Aplikacje często użyje ten parametr podczas ponownego uwierzytelniania już masz wyodrębnionych nazwa użytkownika z poprzedniego logowania przy użyciu `preferred_username` rościć sobie. |
| domain_hint | opcjonalne | Może być jednym z `consumers` lub `organizations`.  Jeśli zawiera, pominie procesu odnajdowania opartej na poczcie e-mail tego użytkownika przez moduł na stronie logowania 2.0, prowadzące do nieco usprawnionym środowisko użytkownika.  Aplikacje często użyje ten parametr podczas ponownego uwierzytelniania po wyodrębnieniu `tid` rościć sobie z id_token.  Jeśli `tid` rościć sobie wartość jest `9188040d-6c67-4c5b-b112-36a304b66dad`, należy użyć `domain_hint=consumers`.  W przeciwnym razie za pomocą `domain_hint=organizations`. |

Na tym etapie użytkownik zostanie poproszony wprowadź swoje poświadczenia i ukończyć uwierzytelniania.  Punkt końcowy 2.0 zapewni również, że użytkownik ma zgodę na uprawnienia określone w `scope` kwerenda parametryczna.  Jeśli użytkownik nie ma zgodę na dowolny z tych uprawnień, zostanie wyświetlone pytanie użytkownikowi wyraża zgodę na wymagane uprawnienia.  Szczegóły dotyczące [uprawnień, zgoda oraz wielu dzierżawy aplikacji znajdują się w tym miejscu](active-directory-v2-scopes.md).

Gdy użytkownik uwierzytelnia i udziela zgody, punkt końcowy 2.0 zwróci odpowiedź dotycząca aplikacji na wskazanej `redirect_uri`, przy użyciu metody określonej w `response_mode` parametru.

#### <a name="successful-response"></a>Pomyślną odpowiedź

Pomyślną odpowiedź przy użyciu `response_mode=fragment` i `response_type=id_token+token` wygląda jak następujących podziałów wierszy dla czytelności:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| access_token | Jeżeli uwzględniane `response_type` zawiera `token`. Token dostępu do tej aplikacji, w tym przypadku dla programu Microsoft Graph.  Token dostępu nie powinny zostać odczytany lub w przeciwnym razie inspekcji, mogą być traktowane jako ciąg jest nieprzezroczysta. |
| token_type | Jeżeli uwzględniane `response_type` zawiera `token`.  Zawsze będzie `Bearer`. |
| expires_in | Jeżeli uwzględniane `response_type` zawiera `token`.  Wskazuje liczbę sekund token jest prawidłowe na potrzeby buforowania. |
| zakres | Jeżeli uwzględniane `response_type` zawiera `token`.  Wskazuje, dla której ma obowiązywać access_token następujące zakresy. |
| id_token | Id_token, tej aplikacji. Za pomocą id_token zweryfikować tożsamość użytkownika i Rozpocznij sesję z danym użytkownikiem.  Więcej informacji na temat id_tokens i ich zawartość jest dołączany do [odwołania do tokenu końcowego 2.0](active-directory-v2-tokens.md).  |
| Województwo | Jeśli parametr Stan znajduje się w wezwaniu na tej samej wartości powinien być wyświetlany w odpowiedzi. Aplikacja należy sprawdzić, czy wartości stanu na żądanie i odpowiedź są identyczne. |


#### <a name="error-response"></a>Odpowiedzi komunikat o błędzie
Odpowiedzi na błędy mogą być również wysyłane do `redirect_uri` , aplikacja może podejmie wobec nich odpowiednio:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kodu błędu może służyć do klasyfikowania typów błędów występujących, która umożliwia reagowanie na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania.  |

## <a name="validate-the-idtoken"></a>Sprawdź poprawność id_token
Po prostu otrzymywanie id_token nie wystarcza do uwierzytelnienia użytkownika; należy sprawdzić poprawność podpisu id_token i sprawdź oświadczeniach tokenu na wymagania aplikacji sieci.  Punkt końcowy 2.0 używa [Tokenów Web JSON (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) i szyfrowania kluczem publicznym do podpisania tokeny i sprawdź, czy są prawidłowe.

Możesz wybrać sprawdzić poprawność `id_token` w kliencie kod, ale wspólnych ćwiczenia jest wysłanie `id_token` na serwerze wewnętrznej bazy danych i wykonać sprawdzanie poprawności.  Po zostały poprawności Podpis id_token, istnieje kilka roszczeń, które trzeba będzie do weryfikacji.  Zobacz [odwołania do tokenu 2.0 —](active-directory-v2-tokens.md) Aby uzyskać więcej informacji, w tym [Sprawdzanie poprawności tokenów](active-directory-v2-tokens.md#validating-tokens) i [Ważne informacje o logowaniu klucz przerzucania](active-directory-v2-tokens.md#validating-tokens).  Zalecamy tokeny korzystające z biblioteki do analizowania i sprawdzanie poprawności — co najmniej jeden dostępne dla większości języków i platformach.
<!--TODO: Improve the information on this-->

Możesz również sprawdzić poprawność dodatkowe roszczenia w zależności od aktualnego scenariusza.  Niektóre typowe reguły sprawdzania poprawności obejmują:

- Zapewnianie użytkownika/organizacji po zarejestrowaniu aplikacji.
- Zapewnianie użytkownik ma pisane z wielkiej litery autoryzacja i uprawnienia
- Zapewnianie wytrzymałości uwierzytelniania wystąpił, takich jak uwierzytelnianie wieloskładnikowe.

Aby uzyskać więcej informacji na oświadczeniach w id_token zobacz [odwołania do tokenu końcowego 2.0](active-directory-v2-tokens.md).

Po całkowicie poprawności id_token można rozpocząć sesję z użytkownikiem i użyć roszczeń w id_token, aby uzyskać informacje o użytkowniku w aplikacji.  Te informacje może służyć do wyświetlania rekordów, zezwolenia itd.

## <a name="get-access-tokens"></a>Uzyskiwanie tokeny dostępu

Teraz, gdy użytkownik został logujesz się do aplikacji pojedynczej strony, możesz uzyskać tokeny dostępu dla połączeń sieci web interfejsy API zabezpieczone Azure AD, takich jak [Microsoft Graph](https://graph.microsoft.io).  Nawet jeśli już odebrane tokenu za pomocą `token` response_type, umożliwiają ta metoda umożliwia uzyskanie tokeny do dodatkowych zasobów bez konieczności przekierować użytkownika, aby zalogować się ponownie.

W procesie OpenID Połącz-OAuth normalny sposób jak to wysyłając żądania do 2.0 `/token` punktu końcowego.  Punkt końcowy 2.0 nie obsługuje jednak żądania CORS, więc nawiązywanie połączeń AJAX gromadzenia i odświeżanie tokeny znajduje się poza pytanie.  Zamiast tego można niejawne przepływu w ukrytych iframe uzyskać nowe tokeny dla innych interfejsów API sieci web: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [AZURE.TIP] Spróbuj kopiowania i wklejania poniżej wezwanie na karcie w przeglądarce! (Pamiętaj zamienić `domain_hint` oraz `login_hint` wartości przy użyciu poprawne wartości dla użytkownika)

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| dzierżawy | Wymagane | `{tenant}` Wartość w polu Ścieżka żądania może być używana w celu kontrolowania, kto może zalogować się do aplikacji.  Są dozwolone wartości `common`, `organizations`, `consumers`i dzierżawa identyfikatory.  Aby uzyskać więcej szczegółów zobacz [podstawy Protocol (protokół)](active-directory-v2-protocols.md#endpoints). |
| client_id | Wymagane | Identyfikator aplikacji przypisania aplikacji portalu rejestracji ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)). |
| response_type | Wymagane | Musi zawierać `id_token` dla połączenia OpenID logowania.  Może również obejmować inne response_types, takich jak `code`. |
| redirect_uri | zalecane | Redirect_uri aplikacji, której uwierzytelniania odpowiedzi można wysyłane i otrzymywane przez aplikację.  Dokładnie musi odpowiadać jeden z redirect_uris, które zarejestrowane w portalu, z wyjątkiem musi być zakodowany w adresie url. |
| zakres | Wymagane | Rozdzielany spacjami zakresów.  Pobieranie tokeny obejmuje wszystkie [zakresy](active-directory-v2-scopes.md) wymaganych dla zasobu zainteresowania.  |
| response_mode | zalecane | Określa metodę, których należy używać wysyłanie uzyskanego wstecz token do aplikacji.  Może być jednym z `query`, `form_post`, lub `fragment`.  |
| Województwo | zalecane | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token.  Może być ciągiem dowolną zawartość, która ma.  Losowo wygenerowanym wartości unikatowe jest zazwyczaj używany służące do zapobiegania powstawaniu atakami fałszowaniu żądanie między witrynami.  Stan służy także do kodowania informacji o stanie użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony lub widoku, w których znajdowały się na. |
| Identyfikator jednorazowy | Wymagane | Wartość zawarte w wezwaniu na wygenerowane za pomocą aplikacji, które zostaną uwzględnione w wyniku id_token jako roszczenia.  Aplikacja może zweryfikować tę wartość na zmniejszeniu atakami token powtarzania.  Wartość jest zazwyczaj te, unikatowy ciąg, który może służyć do identyfikowania początku żądania.  |
| wiersz | Wymagane | Odświeżanie i uzyskiwanie tokeny w ukrytych iframe, należy używać `prompt=none` zapewnienie elementu iframe nie są wysuwane na stronie logowania 2.0 i zwraca natychmiast. |
| login_hint | Wymagane | Odświeżanie i uzyskiwanie tokeny w ukrytych iframe, nazwa użytkownika w wskazówka należy uwzględnić w celu odróżnienia wiele sesji, który użytkownik może korzystać w danym punkcie w czasie. Nazwa użytkownika można wyodrębnić z poprzedniego logowania przy użyciu `preferred_username` rościć sobie. |
| domain_hint | Wymagane | Może być jednym z `consumers` lub `organizations`.  Odświeżanie i uzyskiwanie tokeny w ukrytych iframe, możesz umieścić domain_hint w wezwaniu.  Należy wyodrębnić `tid` rościć sobie z id_token z poprzedniego logowania do określenia wartość, która umożliwia.  Jeśli `tid` rościć sobie wartość jest `9188040d-6c67-4c5b-b112-36a304b66dad`, należy użyć `domain_hint=consumers`.  W przeciwnym razie za pomocą `domain_hint=organizations`. |

Podziękowania dla `prompt=none` parametr, to żądanie zostanie albo powiodła się lub od razu się nie powieść i wrócić do aplikacji.  Pomyślnie odpowiedzi będą wysyłane do aplikacji na wskazanej `redirect_uri`, przy użyciu metody określonej w `response_mode` parametru.

#### <a name="successful-response"></a>Pomyślną odpowiedź
Pomyślną odpowiedź przy użyciu `response_mode=fragment` wygląda jak:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| access_token | Token tej aplikacji. |
| token_type | Zawsze będzie `Bearer`. |
| Województwo | Jeśli parametr Stan znajduje się w wezwaniu na tej samej wartości powinien być wyświetlany w odpowiedzi. Aplikacja należy sprawdzić, czy wartości stanu na żądanie i odpowiedź są identyczne. |
| expires_in | Jak długo trwa token dostępu jest prawidłowy (w sekundach). |
| zakres | Zakresy, które token dostępu dotyczy. |

#### <a name="error-response"></a>Odpowiedzi komunikat o błędzie
Odpowiedzi na błędy mogą być również wysyłane do `redirect_uri` , aplikacja może je prawidłowo obsługiwać.  W przypadku programu `prompt=none`, będzie oczekiwany błąd:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kod błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania.  |

Jeśli zostanie wyświetlony ten błąd w wezwaniu iframe, użytkownik interaktywnie zalogować się ponownie pobrać nowy token.  Istnieje możliwość obsługę w tym przypadku w sposób sens dla aplikacji.

## <a name="refreshing-tokens"></a>Odświeżanie tokenów

Obie `id_token`s i `access_token`s wygaśnie po krótkim czas, więc aplikacji muszą mieć możliwość odświeżania tych tokenów okresowo.  Aby odświeżyć dowolnego typu token, można wykonywać wezwanie na tym samym ukrytych iframe miejsc położonych powyżej, za pomocą `prompt=none` parametr w celu sterowania zachowaniem Azure AD.  Jeśli chcesz otrzymywać nową `id_token`, należy użyć `response_type=id_token` i `scope=openid`, a także `nonce` parametru.


## <a name="send-a-sign-out-request"></a>Wysyłanie Wyloguj żądanie

OpenIdConnect `end_session_endpoint` nie jest obecnie obsługiwane przez punkt końcowy 2.0. Oznacza to, że aplikacji nie można wysłać żądanie do punktu końcowego 2.0, aby zakończyć sesji użytkownika i usunąć pliki cookie, ustaw punkt końcowy 2.0.
Do zalogowania się użytkownika, aplikacji można po prostu zakończenie własnej sesji z danym użytkownikiem i pozostaw sesji użytkownika 2.0 — punkt końcowy w niezmieniony.  Przy następnym użytkownik próbuje zalogować się, przy użyciu konta wymienione w ich aktywnie zalogowany zobaczą stronę "Wybierz konto".
Na tej stronie użytkownika mogą być wylogować się z dowolnym kontem kończenie sesji z punktem końcowym 2.0.

<!--

When you wish to sign the user out of the  app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->