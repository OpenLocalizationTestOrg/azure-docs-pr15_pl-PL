<properties
    pageTitle="Omówienie protokołu .NET Azure AD | Microsoft Azure"
    description="W tym artykule opisano, jak za pomocą wiadomości HTTP autoryzacji dostępu do aplikacji sieci web oraz interfejsy API sieci web w dzierżawie za pomocą usługi Azure Active Directory i OAuth 2.0."
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


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>Autoryzuj dostęp do aplikacji sieci web przy użyciu OAuth 2.0 i usługi Azure Active Directory

Azure Active Directory (Azure AD) przy użyciu OAuth 2.0 umożliwia zezwolić na dostęp do aplikacji sieci web oraz interfejsy API sieci web w dzierżawie usługi Azure AD. Ten przewodnik jest niezależny od języka i opisano, jak wysyłać i odbierać wiadomości HTTP bez przy użyciu jednej z naszych bibliotek Otwórz źródło.

Przepływ kod autoryzacji OAuth 2.0 zostało opisane w [sekcji 4.1 specyfikacji OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-4.1) . Służy do wykonywania uwierzytelniania i autoryzacji w większości typów aplikacji, w tym aplikacji sieci web i oryginalnie zainstalowane aplikacje.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>Przepływ autoryzacji OAuth 2.0

Na wysokim poziomie przepływ całego autoryzacji dla aplikacji wygląda nieco następująco:

![Przepływ uwierzytelniania kod OAuth](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>Żądanie kodu autoryzacji

Przepływ kodu autoryzacji zaczyna się od klienta kierujące użytkownikowi `/authorize` punktu końcowego. W wezwaniu na ten klient wskazuje uprawnienia, które trzeba ją uzyskać od użytkownika. Punkty końcowe OAuth 2.0 można uzyskać na stronie aplikacji w klasycznym Portal Azure, przycisk **Widok punkty końcowe** w szufladzie dołu.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------- |
| dzierżawy | Wymagane | `{tenant}` Wartość w polu Ścieżka żądania może być używana w celu kontrolowania, kto może zalogować się do aplikacji.  Dozwolone wartości są identyfikatorów dzierżawy, na przykład `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` lub `contoso.onmicrosoft.com` lub `common` tokeny niezależny od dzierżawy |
| client_id | Wymagane | Identyfikator aplikacji przypisane do aplikacji podczas rejestrowania z usługą Azure Active Directory. To można znaleźć w portalu zarządzania Azure. Kliknij w **Usłudze Active Directory**, kliknij katalog, kliknij aplikację i następnie wybierz polecenie **Konfiguruj** |
| response_type | Wymagane | Musi zawierać `code` przepływ kodu autoryzacji. |
| redirect_uri | zalecane | Redirect_uri aplikacji, której uwierzytelniania odpowiedzi można wysyłane i otrzymywane przez aplikację.  Dokładnie musi odpowiadać jeden z redirect_uris, które zarejestrowane w portalu, z wyjątkiem musi być zakodowany w adresie url.  W przypadku aplikacji natywne i urządzeń przenośnych, należy używać wartości domyślnej `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode | zalecane | Określa metodę, których należy używać wysyłanie wyniku wstecz token do aplikacji.  Może być `query` lub `form_post`.  |
| Województwo | zalecane | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token. Losowo wygenerowanym wartości unikatowe jest zazwyczaj używany służące do [zapobiegania powstawaniu atakami fałszowaniu żądanie między witrynami](http://tools.ietf.org/html/rfc6749#section-10.12).  Stan służy także do kodowania informacji o stanie użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony lub widoku, w których znajdowały się na. |
| zasób | opcjonalne | Identyfikator URI identyfikator aplikacji sieci Web interfejsu API (zabezpieczonego zasób). Aby znaleźć identyfikator URI identyfikator aplikacji sieci web API, w portalu zarządzania Azure, kliknij pozycję **Usługi Active Directory**, kliknij katalog, kliknij aplikację, a następnie kliknij **Konfiguruj**. |
| wiersz | opcjonalne |  Wskazuje typ interakcji użytkownika, który jest wymagane.<p> Prawidłowe wartości są następujące: <p> *Zaloguj się*: to monitowany o uwierzytelnienie. <p> *wyrażenie zgody*: zgody użytkownika przyznano, ale należy zaktualizować. Użytkownik powinien wyświetlony monit o zgodę. <p> *admin_consent*: administrator powinien zostać wyświetlony monit o zgodę w imieniu wszystkich użytkowników w organizacji |
| login_hint | opcjonalne | Można wstępnie wypełnianie pola Adres nazwa_użytkownika/e-mail stronie logowania dla użytkownika, jeśli znasz jego nazwę użytkownika wyprzedzeniem.  Aplikacje często użyje ten parametr podczas ponownego uwierzytelniania już masz wyodrębnionych nazwa użytkownika z poprzedniego logowania przy użyciu `preferred_username` rościć sobie. |
| domain_hint | opcjonalne | Zawiera wskazówki dotyczące dzierżawy lub domenę, którą użytkownika należy używać do zalogowania się. Wartość argumentu domain_hint jest zarejestrowanych domen dla dzierżawy. Jeśli do katalogu lokalnego Federacji dzierżawy, AAD przekierowuje się na serwerze federacyjnym określoną dzierżawy. |

> [AZURE.NOTE] Jeśli użytkownik jest częścią organizacji, administrator organizacji może wyraża zgodę odrzucić w imieniu użytkownika lub umożliwić użytkownikowi do wyraża zgodę. Użytkownik będzie mieć możliwość zgodę tylko wtedy, gdy administrator pozwala.

Na tym etapie użytkownik zostanie poproszony o wprowadź swoje poświadczenia i wyraża zgodę na wskazany uprawnienia `scope` kwerenda parametryczna. Gdy użytkownik uwierzytelnia i udziela zgody, Azure AD wysyła odpowiedź dotycząca aplikacji u `redirect_uri` adres w wezwaniu.

### <a name="successful-response"></a>Pomyślną odpowiedź

Pomyślną odpowiedź może wyglądać tak:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parametr | Opis |
| -----------------------| --------------- |
| admin_consent | Ma wartość True, jeśli administrator zgodę na monit żądanie zgody.|
| Kod | Kod autoryzacji tej aplikacji. Aplikacja można używać kodu autoryzacji żądania token dostępu dla zasobu docelowego. |
| session_state | Wartość unikatowa, który identyfikuje bieżącej sesji użytkownika. Ta wartość jest identyfikator GUID, ale powinny być traktowane jako wartość nieprzezroczystości, co wartość przekazywana bez badania. |
| Województwo | Jeśli parametr Stan znajduje się w wezwaniu na tej samej wartości powinien być wyświetlany w odpowiedzi. Jest zalecane dla aplikacji, aby zweryfikować, że wartości stanu na żądanie i odpowiedź są identyczne przed użyciem odpowiedź. Dzięki temu wykrywanie [atakami międzywitrynowe żądanie fałszowaniu (CSRF)](https://tools.ietf.org/html/rfc6749#section-10.12) przed klienta.  

### <a name="error-response"></a>Odpowiedzi komunikat o błędzie

Odpowiedzi na błędy mogą być również wysyłane do `redirect_uri` tak, aby aplikacja może podejmie wobec nich prawidłowo.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametr | Opis |
|-----------|-------------|
| Błąd | Wartość kodu błędu zdefiniowane w sekcji 5,2 [Framework autoryzacji 2.0 OAuth](http://tools.ietf.org/html/rfc6749). W następnej tabeli opisano kody błędów, które zwraca Azure AD. |
| error_description | Bardziej szczegółowy opis błędu. Ten komunikat nie ma być przyjazne dla użytkownika końcowego. |
| Województwo | Wartość stanu jest losowo wygenerowanym-ponownie wartość jest wysyłana w wezwaniu na i zwrócony w odpowiedzi na przed atakami fałszowaniu (CSRF) między witrynami wezwanie. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Kody błędów autoryzacji punktu końcowego błędów

W poniższej tabeli opisano różne kody błędów, które mogą zostać zwrócone w `error` parametru odpowiedzi błędu.

| Kod błędu | Opis | Akcja klienta |
|------------|-------------|---------------|
| invalid_request | Błąd protokołu, taki jak brak wymaganego parametru. | Rozwiąż i prześlij żądanie. To jest rozwinięcie błąd jest zwykle otrzymanych podczas testowania początkowej.|
| unauthorized_client | Aby zażądać kod autoryzacji nie jest dozwolona z aplikacją kliencką. | Zwykle dzieje się tak, gdy aplikacja klienta nie jest zarejestrowany w Azure AD lub nie są dodawane do dzierżawy Azure AD użytkownika. Aplikacja może Monituj o instrukcje dotyczące instalowania aplikacji i dodanie go do Azure AD. |
| ACCESS_DENIED | Odmowa zgody właściciela zasobu | Z aplikacją kliencką może powiadomić użytkownika, którego nie można kontynuować, chyba że użytkownik wyraża zgodę. |
| unsupported_response_type | W wezwaniu na serwerze autoryzacji nie obsługuje typ odpowiedzi. | Rozwiąż i prześlij żądanie. To jest rozwinięcie błąd jest zwykle otrzymanych podczas testowania początkowej.|
|server_error | Serwer wystąpił nieoczekiwany błąd. | Ponawianie żądania. Tych błędów może spowodować tymczasowe warunków. Z aplikacją kliencką mogą wyjaśnić użytkownikowi, że odpowiedzi jest opóźnione ukończenia tymczasowy błąd. |
| temporarily_unavailable | Serwer jest tymczasowo zajęta do obsługi żądania. | Ponawianie żądania. Z aplikacją kliencką może wyjaśniono użytkownikowi, że odpowiedzi jest opóźnione ukończenia tymczasowe warunku. |
| invalid_resource |Zasobu docelowego jest nieprawidłowy, ponieważ nie istnieje, nie można go znaleźć Azure AD lub nie jest poprawnie skonfigurowany.| Oznacza to, że zasobu, jeśli istnieje, nie został skonfigurowany w dzierżawie. Aplikacja może Monituj o instrukcje dotyczące instalowania aplikacji i dodanie go do Azure AD. |

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Aby zażądać token dostępu przy użyciu kodu autoryzacji

Teraz, gdy zostało nabyte kod autoryzacji i udzielono uprawnień przez użytkownika mogą zrealizować kod token dostępu do żądanego zasobu przez wysłanie żądania POST do `/token` punkt końcowy:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parametr | | Opis |
| ----------------------- | ------------------------------- | --------------------- |
| dzierżawy | Wymagane |  `{tenant}` Wartość w polu Ścieżka żądania może być używana w celu kontrolowania, kto może zalogować się do aplikacji.  Dozwolone wartości są identyfikatorów dzierżawy, na przykład `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` lub `contoso.onmicrosoft.com` lub `common` tokeny niezależny od dzierżawy |
| client_id | Wymagane | Identyfikator aplikacji przypisane do aplikacji podczas rejestrowania z usługą Azure Active Directory. To można znaleźć w portalu klasycznym Azure. Kliknij w **Usłudze Active Directory**, kliknij katalog, kliknij aplikację i następnie wybierz polecenie **Konfiguruj** |
| grant_type | Wymagane | Musi być `authorization_code` dla przepływu kodu autoryzacji. |
| Kod | Wymagane | `authorization_code` Którego został zakupiony w poprzedniej sekcji   |
| redirect_uri | Wymagane | Taki sam `redirect_uri` wartości, który został użyty do uzyskania `authorization_code`. |
| client_secret | wymagane dla aplikacji sieci web | Hasło aplikacji utworzonej w portalu rejestracji aplikacji dla aplikacji.  Nie należy można użyć w natywnej aplikacji, ponieważ client_secrets nie mogą być w wiarygodny sposób przechowywane na urządzeniach.  Jest to wymagane dla aplikacji sieci web i interfejsów API, które mogą być przechowywane w sieci web `client_secret` bezpieczne po stronie serwera. |
| zasób | wymagane, jeśli określony w wezwaniu na kod autoryzacji jeszcze opcjonalne | Identyfikator URI identyfikator aplikacji sieci Web interfejsu API (zabezpieczonego zasób).
Znajdowanie identyfikatora URI identyfikator aplikacji, w portalu zarządzania Azure, kliknij **Usługi Active Directory**, kliknij katalog, kliknij aplikację, a następnie kliknij przycisk **Konfiguruj**.

### <a name="successful-response"></a>Pomyślną odpowiedź

Azure AD zwraca token dostępu po pomyślnym odpowiedź. Aby zminimalizować sieci połączenia z aplikacją kliencką i jego skojarzony czas oczekiwania, z aplikacją kliencką należy pamięci podręcznej tokeny dostępu dla okres ważności tokenu, którą określono w odpowiedzi OAuth 2.0. Aby określić okres ważności tokenu, użyj jednej `expires_in` lub `expires_on` wartości parametrów.

Jeśli zwraca web zasobów interfejsu API `invalid_token` kod błędu, może to oznaczać, że zasobu wykrył wygasł tokenu. Jeśli godziny zegara klienta i zasobów są różne (nazywane "time skośność"), zasób warto rozważyć token wygasła, zanim token jest usuwane z pamięci podręcznej klienta. W takim przypadku wyczyść tokenu z pamięci podręcznej, nawet jeśli jest nadal w użytkowania obliczeniowe.

Pomyślną odpowiedź może wyglądać tak:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| access_token | Token dostępu wymagane. Aplikacja umożliwia ten token uwierzytelniania zabezpieczone zasobu, takiego jak interfejsu API sieci web. |
| token_type | Wskazuje typ tokenu wartość. Tylko typ, która obsługuje Azure AD jest okaziciela. Aby uzyskać więcej informacji na temat tokeny okaziciela zobacz [Framework autoryzacji OAuth2.0: zastosowania Token okaziciela (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | Jak długo trwa token dostępu jest prawidłowy (w sekundach). |
| expires_on | Czas wygaśnięcia token dostępu. Data jest reprezentowana jako liczbę sekund od 1970-01-01T0:0:0Z UTC czasu wygaśnięcia. Ta wartość jest używana do określenia ważności tokeny pamięci podręcznej. |
| zasób | Identyfikator URI identyfikator aplikacji sieci Web interfejsu API (zabezpieczonego zasób).|
| zakres | Personifikacji uprawnienia do aplikacji klienckiej. Domyślne uprawnienia `user_impersonation`. Właściciel zabezpieczonego zasobu można rejestrować dodatkowe wartości Azure AD.|
| refresh_token |  Token OAuth 2.0 odświeżania. Aplikacja umożliwia uzyskiwanie tokeny dostępu dodatkowe po wygaśnięciu bieżącego tokenu dostępu token ten.  Odświeżanie tokeny są długotrwałe i pozwalają zachować dostęp do zasobów przez dłuższy czas. |
| id_token | Niepodpisany JSON Web Token (JWT). Base64Url mogą aplikacji dekodowanie segmenty ten token na żądanie informacji o użytkownik zalogowany. Aplikacja może pamięci podręcznej wartości i wyświetlać je, ale go nie będą miały od nich autoryzacji lub ograniczenia zabezpieczeń. |

### <a name="jwt-token-claims"></a>Roszczeń Token JWT
Token JWT na wartość `id_token` parametru może zostać odczytany na oświadczeniach następujące czynności:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

`id_token` Parametr zawiera następujące typy oświadczeń. Aby uzyskać więcej informacji o tokeny web JSON zobacz [Specyfikacja wersja robocza JWT IETF](http://go.microsoft.com/fwlink/?LinkId=392344). Aby uzyskać więcej informacji na temat typów token i roszczeń przeczytaj [obsługiwane Token i typy oświadczeń](active-directory-token-and-claims.md)

| Przejmowanie typu | Opis |
|------------|-------------|
| lub | Grupy odbiorców tokenu. Token jest do aplikacji klienta, gdy jest odbiorców `client_id` klienta.
| EXP | Czas wygaśnięcia. Czas wygaśnięcia tokenu. Dla tokenu ważność bieżącej daty/godziny musi być mniejsza niż lub równa argumentowi `exp` wartość. Czas jest reprezentowana przez liczbę sekund od 1 stycznia 1970 (1970-01-01T0:0:0Z) UTC czasu token został wystawiony. |
| family_name | Użytkownika imienia lub nazwiska. Aplikacja może wyświetlać tę wartość. |
| given_name | Imię użytkownika. Aplikacja może wyświetlać tę wartość. |
| IAT | Wygenerowane w czasie. Czas, kiedy JWT został wydany. Czas jest reprezentowana przez liczbę sekund od 1 stycznia 1970 (1970-01-01T0:0:0Z) UTC czasu token został wystawiony. |
| firmy | Służy do identyfikowania wystawcy tokenów |
| NBF | Nie wcześniej niż czas. Czas, kiedy sporządzaniu tokenu. Aby token ma obowiązywać bieżącej daty/godziny musi być większa lub równa wartości Nbf. Czas jest reprezentowana przez liczbę sekund od 1 stycznia 1970 (1970-01-01T0:0:0Z) UTC czasu token został wystawiony. |
| OID | Obiekt identyfikator obiektu użytkownika w Azure AD. |
| Sub | Identyfikator token tematu. To jest trwałe i niezmienne identyfikator użytkownika, który opisuje tokenu. Za pomocą tej wartości w pamięci podręcznej logicznych. |
| TID | Dzierżawa której wystawiony token dzierżawy Azure AD identyfikator (ID). |
| unique_name | Unikatowy identyfikator, które mogą być wyświetlane użytkownikowi. Jest to zazwyczaj głównej nazwy użytkownika (UPN). |
| głównej nazwy użytkownika | Główna nazwa użytkownika użytkownika. |
| Wersja | Wersja. Wersja tokenu JWT, zwykle 1.0. |

### <a name="error-response"></a>Odpowiedzi komunikat o błędzie

Błędy punktu końcowego wydawania token są kody błędów HTTP, ponieważ klient nawiąże połączenie z punktu końcowego wydawania token bezpośrednio. Oprócz kod stanu HTTP punktu końcowego wydawania token Azure AD zwraca JSON dokumentu z obiektów, które dobrze opisują komunikat o błędzie.

Przykładowe odpowiedź błędu może wyglądać tak:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kod błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania.  |
| error_codes | Lista kodów błędów określonych usługi STS, które mogą ułatwić diagnostyki. |
| Sygnatura czasowa | Czas, w którym wystąpił błąd. |
| trace_id | Unikatowy identyfikator żądania, które mogą być pomocne w diagnostyczne.  |
| correlation_id | Unikatowy identyfikator żądania, które mogą być pomocne w diagnostyczne elementów.|

#### <a name="http-status-codes"></a>Kody stanu HTTP

Poniższa tabela zawiera listę kodów stanu HTTP, które zwraca punktu końcowego wydawania token. W niektórych przypadkach kod błędu wystarcza do opisują odpowiedź, ale w przypadku wystąpienia błędów, będzie konieczne analizy towarzyszące dokumentu JSON i sprawdź jego kod błędu.

| Kod HTTP | Opis |
|-----------|-------------|
| 400       | Domyślny kod HTTP. Używane w większości przypadków i zwykle jest to żądanie nieprawidłowo. Rozwiąż i prześlij żądanie. |
| 401       | Uwierzytelnianie nie powiodło się. Na przykład żądanie Brak parametru client_secret.|
| 403       | Autoryzacja nie powiodła się. Na przykład użytkownik nie ma uprawnień dostępu do tego zasobu. |
| 500       | Wystąpił błąd wewnętrzny w punkcie usług. Ponawianie żądania. |

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

## <a name="use-the-access-token-to-access-the-resource"></a>Uzyskiwanie dostępu do zasobu za pomocą token dostępu

Teraz, gdy został pomyślnie pozyskano `access_token`, można używać tokenu w żądania do interfejsów API sieci Web, umieszczając go w `Authorization` nagłówka. Specyfikacja [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) wyjaśniono, jak używać tokeny okaziciela w żądania HTTP dostępu do chronionych zasobów.

### <a name="sample-request"></a>Przykładowe żądanie

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Odpowiedzi komunikat o błędzie

Zasoby zabezpieczone implementujących kody stanu HTTP problem RFC 6750. Jeśli żądanie nie zawiera poświadczeń uwierzytelniania lub brak tokenu, odpowiedź zawiera `WWW-Authenticate` nagłówka. W przypadku niepowodzenia żądanie, serwer zasobu odpowiada z kodem stanu HTTP i kod błędu.

Oto przykład niepowodzenie odpowiedzi podczas żądanie klienta nie zawiera token okaziciela:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Błąd parametrów

| Parametr | Opis |
|-----------|-------------|
| authorization_uri | Identyfikator URI (fizycznie punkt końcowy) na serwerze autoryzacji. Ta wartość również jest używany jako klucz wyszukiwania Aby uzyskać więcej informacji na temat serwera z punkt końcowy odnajdowania. <p><p> Klienta należy sprawdzić, czy na serwerze autoryzacji jest zaufanych. Zasób jest chroniony Azure AD, wystarczy sprawdzić, czy adres URL zaczyna się od https://login.windows.net lub innej nazwy hosta, która obsługuje Azure AD. Zasób specyficzne dla dzierżawy zawsze powinna zwrócić autoryzacji specyficzne dla dzierżawy identyfikatora URI. |
| Błąd | Wartość kodu błędu zdefiniowane w sekcji 5,2 [Framework autoryzacji 2.0 OAuth](http://tools.ietf.org/html/rfc6749).|
| error_description | Bardziej szczegółowy opis błędu. Ten komunikat nie ma być przyjazne dla użytkownika końcowego.|
| Identyfikator_zasobu_2 | Zwraca unikatowy identyfikator zasobu. Z aplikacją kliencką może użyć tego identyfikatora jako wartość `resource` parametru w przypadku żądania tokenu dla zasobu. <p><p> Ważne do aplikacji klienckiej sprawdzić, ta wartość jest, w przeciwnym razie niebezpieczną usługą może być możliwe wywołać atakiem **umożliwia podniesienie uprawnień** <p><p> Sprawdź, czy jest zalecana strategia służące do zapobiegania powstawaniu atakiem `resource_id` odpowiada podstawy adres URL interfejsu API sieci web, który jest dostępny. Jeśli https://service.contoso.com/data jest dostępny, na przykład `resource_id` może być htttps://service.contoso.com/. Aplikacja klienta musi odrzucić `resource_id` nie zaczynające się od podstawowy adres URL nie istnieje zaufanego innym sposobem Sprawdź identyfikator. |

#### <a name="bearer-scheme-error-codes"></a>Kody błędów systemu okaziciela

Specyfikacja RFC 6750 definiuje następujące błędy dla zasobów, które za pomocą za pomocą uwierzytelniania "www" Nagłówek i schemat okaziciela w odpowiedzi.

| Kod stanu HTTP | Kod błędu | Opis | Akcja klienta |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | Żądanie nie jest poprawny. Na przykład może być brak parametru lub przy użyciu tego samego parametru dwa razy. | Naprawianie błędu, a następnie ponów żądanie. Ten typ błędu powinna zostać wykonana tylko w czasie projektowania i wykrywania podczas testowania początkowej. |
| 401 | invalid_token   | Token dostępu został utracony, nieprawidłowe lub został odwołany. Wartość parametru error_description udostępnia dodatkowe informacje. |  Żądanie nowego tokenu z serwera autoryzacji. Jeśli nie powiedzie się nowy token wystąpił nieoczekiwany błąd. Wyślij komunikat o błędzie do użytkownika i spróbuj ponownie po opóźnienia losowe. |
| 403 | insufficient_scope | Token dostępu nie zawiera personifikacji uprawnienia wymagane do uzyskiwania dostępu do zasobu. | Wysłać żądanie nowego autoryzacji do punktu końcowego autoryzacji. Jeśli odpowiedź zawiera parametr zakresu, użyj wartości z zakresu w wezwaniu do zasobu. |
| 403 | insufficient_access | Temat tokenu nie ma uprawnień wymaganych do uzyskania dostępu do zasobu. | Monituje użytkownika za pomocą innego konta albo zażądaj uprawnień do określonego zasobu. |

## <a name="refreshing-the-access-tokens"></a>Odświeżanie tokeny dostępu

Tokeny dostępu są krótkotrwałe i muszą zostać odświeżone po ich wygaśnięciu, aby kontynuować, uzyskiwanie dostępu do zasobów. Można odświeżać `access_token` przesyłając innego `POST` żądanie `/token` punktu końcowego, ale ten czas, dostarczając `refresh_token` zamiast `code`.

Odświeżanie tokeny nie mają określonego istnienia. Zazwyczaj okresów tokeny odświeżania są stosunkowo długich. Jednak w niektórych przypadkach tokeny odświeżania wygaśnie, są odwołany lub brak odpowiednich uprawnień do odpowiedniej akcji. Aplikacja należy się spodziewać i obsługi błędów prawidłowo zwracane przez końcowego wydawania token.

Po otrzymaniu odpowiedzi z błędem token odświeżania odrzucenie bieżącej token odświeżania i żądanie nowy kod autoryzacji lub token dostępu. W szczególności kiedy za pomocą odświeżania token w procesie udzielanie kod autoryzacji Jeśli otrzymasz odpowiedź z `interaction_required` lub `invalid_grant` kody błędów, Odrzuć token odświeżania i poproś o nowy kod autoryzacji.

Próbki żądania do punktu końcowego **specyficzne dla dzierżawy** (możesz także użyć **typowych** punktu końcowego) uzyskanie nowego dostępu token przy użyciu tokenu odświeżania wygląda następująco:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Parametr | Opis |
|-----------|-------------|
| access_token | Nowy token dostępu zażądano.|
| expires_in   | Pozostały czas użytkowania tokenu, w sekundach. Typowa wartość to 3600 (jedna godzina). |
| expires_on   | Data i czas wygaśnięcia tokenu. Data jest reprezentowana jako liczbę sekund od 1970-01-01T0:0:0Z UTC czasu wygaśnięcia. |
| refresh_token | Nowy refresh_token OAuth 2.0, których można zażądać nowego tokeny dostępu po wygaśnięciu w tej odpowiedzi. |
| zasób     | Identyfikuje zabezpieczonego zasób, którego token dostępu może być używana do programu access. |
| zakres        | Personifikacji uprawnienia do natywnej aplikacji. Domyślne uprawnienia jest **user_impersonation**. Wartości alternatywne można rejestrować Azure AD właściciela zasobu docelowego. |
| token_type   | Typ tokenu. Jedyna obsługiwana wartość to **okaziciela**. |

### <a name="successful-response"></a>Pomyślną odpowiedź

Pomyślną odpowiedź token będzie wyglądać:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>Odpowiedzi komunikat o błędzie

Przykładowe odpowiedź błędu może wyglądać tak:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kod błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania.  |
| error_codes | Lista kodów błędów określonych usługi STS, które mogą ułatwić diagnostyki. |
| Sygnatura czasowa | Czas, w którym wystąpił błąd. |
| trace_id | Unikatowy identyfikator żądania, które mogą być pomocne w diagnostyczne.  |
| correlation_id | Unikatowy identyfikator żądania, które mogą być pomocne w diagnostyczne elementów.|

Opis akcji zalecane klienta i kody błędów zobacz [kody błędów pod kątem błędów token punktu końcowego](#error-codes-for-token-endpoint-errors).
