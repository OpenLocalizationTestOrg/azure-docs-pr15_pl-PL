<properties
    pageTitle="Omówienie protokołu .NET Azure AD | Microsoft Azure"
    description="W tym artykule opisano, jak za pomocą wiadomości HTTP autoryzacji dostępu do aplikacji sieci web oraz interfejsy API sieci web w dzierżawie za pomocą usługi Azure Active Directory i łączenie OpenID."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Autoryzuj dostęp do aplikacji sieci web przy użyciu połączenia OpenID i usługi Azure Active Directory

[Łączenie OpenID](http://openid.net/specs/openid-connect-core-1_0.html) to warstwa prosty tożsamości wbudowane na wierzchu protokołu OAuth 2.0. OAuth 2.0 definiuje mechanizmy do uzyskiwanie dostępu do chronionych zasobów za pomocą **tokeny dostępu** , ale nie definiują standardowe metody o podanie informacji o tożsamości. Łączenie OpenID uwierzytelnianie jest realizowanie jako rozszerzenie proces autoryzacji OAuth 2.0, dostarczając informacji o użytkownika końcowego w postaci `id_token` która sprawdza tożsamość użytkownika, a także udostępnia podstawowe informacje o profilu użytkownika.

Łączenie OpenID jest naszych rekomendacji, jeśli tworzysz aplikację sieci web znajdującej się na serwerze i uzyskać do nich dostęp za pomocą przeglądarki sieci.

## <a name="authentication-flow-using-openid-connect"></a>Przepływ uwierzytelniania przy użyciu funkcji OpenID połączenie

Najprostszym przepływ logowania zawiera następujące czynności — każdy z nich jest opisane szczegółowo poniżej.

![Łączenie OpenId przepływ uwierzytelniania](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## <a name="send-the-sign-in-request"></a>Wyślij żądanie logowania

Gdy aplikacja sieci web wymaga do uwierzytelnienia użytkownika, należy skierować użytkownika `/authorize` punktu końcowego. Wniosek jest podobne do pierwszego gałęzi [OAuth 2.0 autoryzacji kod przepływ](active-directory-protocols-oauth-code.md), z kilku ważnych różnice:

- Żądanie musi zawierać zakres `openid` w `scope` parametru.
- `response_type` Parametr musi zawierać `id_token`.
- Żądanie musi zawierać `nonce` parametru.

Aby przykładowe żądanie będzie miała następującą postać:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| dzierżawy | Wymagane | `{tenant}` Wartość w polu Ścieżka żądania może być używana w celu kontrolowania, kto może zalogować się do aplikacji.  Dozwolone wartości są identyfikatorów dzierżawy, na przykład `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` lub `contoso.onmicrosoft.com` lub `common` tokeny niezależny od dzierżawy |
| client_id | Wymagane | Identyfikator aplikacji przypisane do aplikacji podczas rejestrowania z usługą Azure Active Directory. To można znaleźć w portalu klasyczny Azure. Kliknij w **Usłudze Active Directory**, kliknij katalog, kliknij na pasku aplikacji i następnie wybierz polecenie **Konfiguruj** |
| response_type | Wymagane | Musi zawierać `id_token` dla połączenia OpenID logowania.  Może również obejmować inne response_types, takich jak `code`. |
| zakres | Wymagane | Rozdzielany spacjami zakresów.  Dla OpenID połączyć, musi zawierać zakres `openid`, które przekłada się na uprawnienia "Zalogowanie użytkownika" w zgody interfejsu użytkownika.  Można także dodać inne zakresy w tym żądaniu żąda zgody. |
| Identyfikator jednorazowy | Wymagane | Wartość zawarte w wezwaniu na wygenerowane za pomocą aplikacji, które zostaną uwzględnione w celu utworzenia `id_token` jako roszczenia.  Aplikacja może zweryfikować tę wartość na zmniejszeniu atakami token powtarzania.  Wartość jest zazwyczaj te unikatowy ciąg lub identyfikator GUID, który może być używany do identyfikowania początku żądania.  |
| redirect_uri | zalecane | Redirect_uri aplikacji, której uwierzytelniania odpowiedzi można wysyłane i otrzymywane przez aplikację.  Dokładnie musi odpowiadać jeden z redirect_uris, które zarejestrowane w portalu, z wyjątkiem musi być zakodowany w adresie url. |
| response_mode | zalecane | Określa metodę, które powinny być używane do wysyłania wyniku authorization_code powrót do aplikacji.  Obsługiwane są następujące wartości `form_post` dla *Publikowanie formularza HTTP* lub `fragment` dla *fragment adresu URL*.  Dla aplikacji sieci web zaleca się używanie `response_mode=form_post` zapewnienie transferu najbezpieczniejsza tokenów do aplikacji.  
| Województwo | zalecane | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token.  Może być ciągiem dowolną zawartość, która ma.  Losowo wygenerowanym wartości unikatowe jest zazwyczaj używany służące do [zapobiegania powstawaniu atakami fałszowaniu żądanie między witrynami](http://tools.ietf.org/html/rfc6749#section-10.12).  Stan służy także do kodowania informacji o stanie użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony lub widoku, w których znajdowały się na. |
| wiersz | opcjonalne | Wskazuje typ interakcji użytkownika, który jest wymagane.  Jedyne prawidłowe wartości w tej chwili to "logowania", "Brak" i "zgoda".  `prompt=login`będzie wymusić na użytkowniku wprowadź swoje poświadczenia na wniosku, Negacja logowania jednokrotnego na.  `prompt=none`odwrotny - go będziesz mieć pewność, że użytkownik nie jest wyświetlany wszystkie interaktywne monity jakiejkolwiek.  Jeśli żądanie nie może się zakończyć w trybie cichym za pośrednictwem logowania jednokrotnego na, punkt końcowy zwróci błąd.  `prompt=consent`wywoła okno dialogowe zgody OAuth, gdy użytkownik zaloguje, użytkownikowi uprawnienia do aplikacji. |
| login_hint | opcjonalne | Można wstępnie wypełnianie pola Adres nazwa_użytkownika/e-mail stronie logowania dla użytkownika, jeśli znasz jego nazwę użytkownika wyprzedzeniem.  Aplikacje często użyje ten parametr podczas ponowne uwierzytelnianie, masz już wyodrębnionych nazwa użytkownika z poprzedniego logowania przy użyciu `preferred_username` rościć sobie. |


Na tym etapie użytkownik zostanie poproszony wprowadź swoje poświadczenia i ukończyć uwierzytelniania.

### <a name="sample-response"></a>Przykładowe odpowiedzi

Odpowiedź przykładowy, gdy użytkownik uwierzytelniony, może wyglądać tak:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| id_token | `id_token` Tej aplikacji. Możesz użyć `id_token` zweryfikować tożsamość użytkownika i Rozpocznij sesję z danym użytkownikiem.  |
| Województwo | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token. Losowo wygenerowanym wartości unikatowe jest zazwyczaj używany służące do [zapobiegania powstawaniu atakami fałszowaniu żądanie między witrynami](http://tools.ietf.org/html/rfc6749#section-10.12).  Stan służy także do kodowania informacji o stanie użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony lub widoku, w których znajdowały się na. |

### <a name="error-response"></a>Odpowiedzi komunikat o błędzie
Odpowiedzi na błędy mogą być również wysyłane do `redirect_uri` , aplikacja może podejmie wobec nich odpowiednio:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kod błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Kody błędów autoryzacji punktu końcowego błędów

W poniższej tabeli opisano różne kody błędów, które mogą zostać zwrócone w `error` parametru odpowiedzi błędu.

| Kod błędu | Opis | Akcja klienta |
|------------|-------------|---------------|
| invalid_request | Błąd protokołu, taki jak brak wymaganego parametru. | Rozwiąż i prześlij żądanie. To jest rozwinięcie błąd jest zwykle otrzymanych podczas testowania początkowych.|
| unauthorized_client | Aby zażądać kod autoryzacji nie jest dozwolona z aplikacją kliencką. | Zwykle dzieje się tak, gdy aplikacja klienta nie jest zarejestrowany w Azure AD lub nie są dodawane do dzierżawy Azure AD użytkownika. Aplikacja może Monituj o instrukcje dotyczące instalowania aplikacji i dodanie go do Azure AD. |
| ACCESS_DENIED | Odmowa zgody właściciela zasobu | Z aplikacją kliencką może powiadomić użytkownika, którego nie można kontynuować, chyba że użytkownik wyraża zgodę. |
| unsupported_response_type | W wezwaniu na serwerze autoryzacji nie obsługuje typu odpowiedzi. | Rozwiąż i prześlij żądanie. To jest rozwinięcie błąd jest zwykle otrzymanych podczas testowania początkowych.|
|server_error | Serwer napotkał nieoczekiwany błąd. | Ponawianie żądania. Tych błędów może spowodować tymczasowe warunków. Z aplikacją kliencką może wyjaśniono użytkownikowi, że odpowiedzi jest opóźnione ukończenia tymczasowy błąd. |
| temporarily_unavailable | Serwer jest tymczasowo zajęta do obsługi żądania. | Ponawianie żądania. Z aplikacją kliencką może wyjaśnienia użytkownikowi, że odpowiedzi jest opóźnione ukończenia tymczasowe warunku. |
| invalid_resource |Zasobu docelowego jest nieprawidłowy, ponieważ nie istnieje, nie można go znaleźć Azure AD lub nie jest poprawnie skonfigurowany.| Oznacza to, że zasobu, jeśli istnieje, nie został skonfigurowany w dzierżawie. Aplikacja może Monituj o instrukcje dotyczące instalowania aplikacji i dodanie go do Azure AD. |

## <a name="validate-the-idtoken"></a>Sprawdź poprawność id_token

Po prostu otrzymywania `id_token` nie wystarcza do uwierzytelnienia użytkownika; należy sprawdzić poprawność podpisu i sprawdź roszczeń w `id_token` na wymagania dotyczące usługi aplikacji. Punkt końcowy Azure AD używa tokenów Web JSON (JWTs) i szyfrowania kluczem publicznym do podpisania tokeny i sprawdź, czy są prawidłowe.

Możesz wybrać sprawdzić poprawność `id_token` w kliencie kod, ale wspólnych ćwiczenia jest wysłanie `id_token` na serwerze wewnętrznej bazy danych i wykonać sprawdzanie poprawności. Po zostały poprawności Podpis `id_token`, istnieje kilka oświadczeń trzeba będzie do weryfikacji.

Możesz również sprawdzić poprawność dodatkowe roszczenia w zależności od aktualnego scenariusza. Niektóre typowe reguły sprawdzania poprawności obejmują:

- Zapewnianie użytkownika/organizacji po zarejestrowaniu aplikacji.
- Zapewnianie użytkownik ma pisane z wielkiej litery autoryzacja i uprawnienia
- Zapewnianie wytrzymałości uwierzytelniania wystąpił, takich jak uwierzytelnianie wieloskładnikowe.

Gdy całkowicie sprawdzana poprawność `id_token`, można rozpocząć sesję z użytkownikiem i używać roszczeń w `id_token` Aby uzyskać informacje o użytkowniku w aplikacji. Te informacje może służyć do wyświetlania, rekordów, zezwoleń itd. Aby uzyskać więcej informacji na temat typów token i roszczeń przeczytaj [obsługiwane Token i typy oświadczeń](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Wysyłanie Wyloguj żądanie

Jeżeli chcesz zarejestrować użytkownika się z aplikacji, nie wystarczy je wyczyścić pliki cookie Twojej aplikacji lub w inny sposób end sesji z danym użytkownikiem.  Należy również przekierować użytkownikowi `end_session_endpoint` do zalogowania się.  Jeśli nie możesz to zrobić, użytkownik będzie mógł ponownie uwierzytelniania aplikacji bez wprowadzania poświadczeń ponownie, ponieważ będą one miały prawidłowy jednej logowania jednokrotnego sesji z punktem końcowym Azure AD.

Po prostu można przekierowywać użytkownikowi `end_session_endpoint` wymienione w dokumencie metadanych OpenID połączenia:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parametr | | Opis |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | zalecane | Adres URL, którego przekierowywany użytkownik aby po pomyślnym Wyloguj.  Jeśli nie jest dostępny, zostanie wyświetlony komunikat ogólny.  |

## <a name="token-acquisition"></a>Acquisition tokenu

Wiele aplikacji sieci web trzeba nie tylko zalogować użytkownika, ale także uzyskać dostęp do usługi sieci web w imieniu tego użytkownika przy użyciu OAuth. W tym scenariuszu łączy łączenie OpenID uwierzytelniania użytkownika podczas pobierania jednocześnie `authorization_code` można uzyskać `access_tokens` za pomocą przepływu kod autoryzacji OAuth.

## <a name="get-access-tokens"></a>Uzyskiwanie tokeny dostępu

Uzyskanie tokeny dostępu, należy zmodyfikować żądania logowania z góry:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e      // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                   
&state=12345                                         // Any value, provided by your app
&nonce=678910                                        // Any value, provided by your app
```

W tym zakresów uprawnień w wezwaniu i używając `response_type=code+id_token`, `authorize` punktu końcowego będziesz mieć pewność, że użytkownik ma zgodę na uprawnienia określone w `scope` kwerendy parametrycznej i zwracają aplikacji kod autoryzacji do wymiany dla token dostępu.

### <a name="successful-response"></a>Pomyślną odpowiedź

Pomyślną odpowiedź przy użyciu `response_mode=form_post` wygląda jak:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| id_token | `id_token` Tej aplikacji. Możesz użyć `id_token` zweryfikować tożsamość użytkownika i Rozpocznij sesję z danym użytkownikiem. |
| Kod | Authorization_code, tej aplikacji. Aplikacja umożliwia kodu autoryzacji żądanie token dostępu dla zasobu docelowego. Authorization_codes są bardzo krótko, zwykle wygasają po około 10 minut. |
| Województwo | Jeśli parametr Stan znajduje się w wezwaniu na tej samej wartości powinien być wyświetlany w odpowiedzi. Aplikacja należy sprawdzić, czy wartości stanu na żądanie i odpowiedź są identyczne. |

### <a name="error-response"></a>Odpowiedzi komunikat o błędzie

Odpowiedzi na błędy mogą być również wysyłane do `redirect_uri` , aplikacja może podejmie wobec nich odpowiednio:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kod błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania.  |

Opis możliwych kodów błędów i ich działania zalecane klienta zobacz [kody błędów autoryzacji punktu końcowego błędów](#error-codes-for-authorization-endpoint-errors).

Po zostały pobrano autoryzacji `code` i `id_token`, możesz zalogować się użytkownika i uzyskać tokeny dostępu w imieniu tej osoby.  Do zalogowania użytkownika w, należy sprawdzić poprawność `id_token` dokładnie tak, jak opisano powyżej. Aby uzyskać tokeny dostępu, można wykonać kroki opisane w naszą [dokumentację protokołu OAuth](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token).
